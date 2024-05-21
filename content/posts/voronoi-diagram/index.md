---
title: "Voronoi Diagram"
date: 2024-05-15
draft: false
summary: ""
tags: ["Explanation", "Voronoi", "Diagram", "Algorithm", "Image", "Computational Geometry"]
---

# Voronoi Diagram Explanation

Voronoi diagrams, also known as Dirichlet tessellation or Thiessen polygons, are everywhere in nature. You’ve likely encountered them thousands of times, but perhaps didn’t know what they were called. Voronoi diagrams are simple, yet they have incredible properties that have applications in fields ranging from cartography, biology, computer science, statistics, archaeology, all the way to architecture and arts.

First, it should be noted that for any positive integer n, there are n-dimensional Voronoi diagrams, but for now we will only be dealing with two-dimensional Voronoi diagrams. The Voronoi diagram of a set of “sites” or “generators” (points) is a collection of regions that divide up the plane. Each region corresponds to one of the sites or generators, and all of the points in one region are closer to the corresponding site than to any other site. Where there is not one closest point, there is a boundary.

As an analogy imagine a Voronoi diagram in R^2 to contain a series of islands(our generator points). Suppose that each of these islands has a boat, with each boat capable of going the same speed. Let every point in R that can be reached from the boat from island x before any other boat can be associated with island x. The region of points associated with island x is called a Voronoi Diagram.

The basic idea of Voronoi Diagram has many applications in fields both within and outside the math world. Voronoi Diagrams can be used both within and outside the math world. Voronoi diagrams can be used as both a method of solving problems or as a model for examples that already exist. They are very useful in Computational Geometry, particularly for representation or quantization problems, and are used in the field of robotics for creating a protocol for avoiding detected obstacles. For modeling natural occurences, they are helpful in the studies of plant competition(echology & forestry), territories of animals(zoology) and neolithic clans and tribes(anthropology and archaelogy), and patterns of urban settelments(geography).

## Voronoi Diagram Definition

Suppose you have n points scattered on a plane, the Voronoi diagram of those points subdivides the plane in exactly n cells enclosing the portion of the plane that is the closest to each point. This produces a tessellation that completely covers the plane. In the illustration below, I plotted 100 random points and their corresponding Voronoi diagram. As you can see, every point is enclosed in a cell, whose boundaries are equidistant between two or more points. In other words, the area enclosed in the cell is closer to the point in the cell than to any other point.

## Voronoi Diagram's History

Voronoi diagrams were considered as early at 1644 by René Descartes and were used by Dirichlet (1850) in the investigation of positive quadratic forms. They were also studied by Voronoi (1907), who extended the investigation of Voronoi diagrams to higher dimensions. They find widespread applications in areas such as computer graphics, epidemiology, geophysics, and meteorology. A particularly notable use of a Voronoi diagram was the analysis of the 1854 cholera epidemic in London, in which physician John Snow determined a strong correlation of deaths with proximity to a particular (and infected) water pump on Broad Street (Snow 1854, Snow 1855). In his analysis, Snow constructed a map on which he drew a line labeled "Boundary of equal distance between Broad Street Pump and other Pumps." This line essentially indicated the Broad Street Pump's Voronoi cell (Austin 2006). However, for an analysis highlighting some of the oversimplifications and misattributions in this folklore history account of the events surrounding Snow and the London cholera incident, see Field (2020).

## In Nature

Voronoi diagram patterns are common in nature. From microscopic cells in onion skins, to the shell of jackfruits and the coat of giraffes, these patterns are everywhere.

A reason for their omnipresence is that they form efficient shapes. As we mentioned earlier, a Voronoi diagram completely tessellates the plane. All space is used. This is very convenient if you are trying to squeeze as much as possible in a limited space — such as in muscle fibers or bee hives. Voronoi diagrams are also a spontaneous pattern whenever something is growing at a uniform growth rate from separate points as in the illustration below. For instance, this explains why giraffes exhibit such a pattern. Giraffe embryos have a scattered distribution of melanin-secreting cells, which is responsible for the dark pigmentation of the giraffe’s spots. Over the course of the gestation these cells release melanin — hence spots radiate outward. A study from researchers Marcelo Walter, Alan Fournier and Menevaux also explores this concept of using Voronoi diagrams to model computer rendering of spots on animal coats.

## In architecture & art

Perhaps because of their spontaneous, natural look, or simply because of their mesmerizing randomness, Voronoi patterns have intentionally been implemented in human-made structures. An architectural example is the “Water cube,” which was built to house water sports during the 2008 Beijing Olympics. It features Voronoi diagrams on its ceiling and façades. The Voronoi diagrams were chosen because they recall bubbles . This analogy is clear at night, when the entire façade is illuminated in blue and comes alive.

But appreciation for the Voronoi pattern is surely older than this building in China. Guan and Ge ware from the Song dynasty have a distinctive crackled glaze. Ceramics can easily crack during the cooling process, however the crackles from the Guan and Ge ware are different because they are intentional. They were sought after because of their aesthetic qualities. Thanks to the Voronoi-like patterns on their surface, each piece is unique. To date, they are one of the most imitated styles of porcelain.

Voronoi diagrams are also common in graphic arts for creating “abstract” patterns. I think they make excellent background images. For example, I created the thumbnail of this post by generating random points and constructing a Voronoi diagram. Then, I coloured each cell based on the distance of its point from a randomly selected spot in the box. Endless abstract backgrounds images could be generated this way.

## Voronoi Diagram & Delaunay Triangulation

The Delaunay triangulation and Voronoi diagram in R^2 are dual to each other in the graph theoretical sense.
