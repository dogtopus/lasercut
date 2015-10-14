# <lasercut.scad>
Module for openscad, allowing 3d models to be created from 2d lasercut parts

## Basic usage

To prepare a simple rectangle use lasercutoutSquare()
```
include <lasercut.scad>; 

thickness = 3.1;
x = 50;
y = 100;
lasercutoutSquare(thickness=thickness, x=x, y=y);
```
![alt tag](https://raw.githubusercontent.com/bmsleight/lasercut/master/readme/example-001.png)

More complex path using the lasercutout()
```
include <lasercut.scad>; 

thickness = 3.1;
x = 50;
y = 100;
points = [[0,0], [x,0], [x,y], [x/2,y], [x/2,y/2], [0,y/2], [0,0]];
lasercutout(thickness=thickness, points = points);
```
![alt tag](https://raw.githubusercontent.com/bmsleight/lasercut/master/readme/example-002.png)

## Simple Tab
```
include <lasercut.scad>; 

thickness = 3.1;
x = 50;
y = 100;
lasercutoutSquare(thickness=thickness, x=x, y=y,
    simple_tabs=[
            [UP, x/2, y],
            [DOWN, x/2, 0]
        ]
    );
    
translate([0,y+20,-thickness]) rotate([90,0,0]) 
    lasercutoutSquare(thickness=thickness, x=x, y=y,
        simple_tab_holes=[
                [MID, x/2-thickness/2, thickness]
            ]
    );
```
![alt tag](https://raw.githubusercontent.com/bmsleight/lasercut/master/readme/example-003.png)

## Captive Nuts
Great for hold laser-cut sheets together.

```
include <lasercut.scad>; 

thickness = 3.1;
x = 50;
y = 100;
lasercutoutSquare(thickness=thickness, x=x, y=y,
    simple_tabs=[
            [UP, x/2, y],
            [DOWN, x/2, 0]
        ],
    captive_nuts=[
            [LEFT, 0, y/2],
            [RIGHT, x, y/2],
            
            ]
);

translate([0,y+20,-thickness]) rotate([90,0,0]) 
    lasercutoutSquare(thickness=thickness, x=x, y=y,
        simple_tab_holes=[
                [MID, x/2-thickness/2, thickness]
            ]
    );

translate([-20,0,0]) rotate([90,0,90]) 
    lasercutoutSquare(thickness=thickness, x=y, y=x,
        captive_nut_holes=[
                [DOWN, y/2, 0]
            ]
    );
```
![alt tag](https://raw.githubusercontent.com/bmsleight/lasercut/master/readme/example-004.png)

##  Box

With four, five or six side. Finger joints - the correct alignment to give a flat edge regardless of numebr of sides.

```
include <lasercut.scad>; 

thickness = 3.1;
x = 50;
y = 50;
z = 75; 


for (sides =[4:6])
{
    color("Gold",0.75) translate([100*(sides-4),0,0]) 
        lasercutoutBox(thickness = thickness, x=x, y=y, z=z, sides=sides);
}

```
![alt tag](https://raw.githubusercontent.com/bmsleight/lasercut/master/readme/example-005.png)

##  Finger Joints

Simple finger joints, (as automatically used in the box above). Parameters are direction, startup tap, even number) so for example [UP, 1, 4] - UP direction, starting with a tab not a gap, four figners. 
```
include <lasercut.scad>; 

thickness = 3.1;
x = 50;
y = 100;
lasercutoutSquare(thickness=thickness, x=x, y=y,
    finger_joints=[
            [UP, 1, 4],
            [DOWN, 1, 4]
        ]
    );

translate([0,y+20,thickness]) rotate([90,0,0]) 
    lasercutoutSquare(thickness=thickness, x=x, y=y,
        finger_joints=[
                [UP, 1, 4],
                [DOWN, 1, 4]
            ]
    );
  
```
![alt tag](https://raw.githubusercontent.com/bmsleight/lasercut/master/readme/example-006.png)

## Add or remove circles, make a slit or cut-out a rectangle
Circles are [radius, x, y]
Slits are [direction,x,y,length of slit]
Cutouts are [x, y, width of cutout, height of cutout]

```
include <lasercut.scad>; 

thickness = 3.1;
x = 50;
y = 100;
r = thickness*2;
lasercutoutSquare(thickness=thickness, x=x, y=y,
    finger_joints=[
            [UP, 1, 4],
            [DOWN, 1, 4]
        ],
    circles_add = [
            [r, x+thickness, y/5],
        ],
    circles_remove = [
            [r, x/2, y/2],
            [1.5, x/2, y*2/3], // Screw-hole
        ],
    slits = [
            [RIGHT,x,y/2,10]
        ],
    cutouts = [
            [x/6, y/6, thickness*5, thickness*2]
        ]
    );
    
```
![alt tag](https://raw.githubusercontent.com/bmsleight/lasercut/master/readme/example-007.png)
    

