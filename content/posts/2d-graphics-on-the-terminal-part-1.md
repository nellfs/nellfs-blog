+++
title = 'Drawing Graphics in the Terminal Using Go (Part 1)'
date = '2025-12-28T14:41:00-03:00'
draft = false
+++

In this series of tutorials, we will focus on the basics of drawing graphics in the terminal. We’ll use Go, but the concepts can be transferred to almost any other language. We’ll start by creating a simple **Canvas** and drawing a background on it. Then, we’ll explore how to draw multiple characters on the Canvas to create animations or more complex graphics.

This is a very basic tutorial, but with these core concepts you can build things like games, text-based interfaces, data visualizations, and even character based 3D graphics in the terminal.

I hope that by the end of this series you’ll be able to create your own terminal-based applications and also understand how libraries like [Charm](https://github.com/charmbracelet) work under the hood :).

{{< video webm="/videos/happy-2026.webm" mp4="/videos/happy-2026.mp4" >}}

---

## The Canvas

First of all, we need to understand *what* we are drawing on. A **Canvas** is simply a place to draw graphics. In our case, it’s a grid of characters. You can think of it as a two-dimensional array where each cell holds a character.

**Here is a canvas:**

```
ㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤ
ㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤ
ㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤ
ㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤ
ㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤㅤ
```

Sorry, this is an empty canvas, but we’ll fill it with characters soon!  
For the sake of clarity, let’s fill this canvas with dots (`.`).

**Visible Canvas:**

```bash
.......... // 10 dots long
..........
..........
..........
..........
// 5 dots high
```

We can say this canvas is a 10x5 canvas, where the width is 10 and the height is 5

**Dimensions:**

```Go
Width: 10
Height: 5
```

We can represent this in Go as a canvas struct:

```Go
type canvas struct {
	Width      int
	Height     int
	Background rune // out dots (`.`)
}
```

Now we can create a simple constructor function to make sure all fields are set correctly:

```Go
func NewCanvas(width, height int, background rune) *canvas {
	return &canvas{
		Width:      width,
		Height:     height,
		Background: background,
	}
}
```

### Creating the Canvas

```Go
package main

import "github.com/nellfs/go2d/internal/screen"
// I've created the screen package to hold the canvas.
// You can organize the code as you want.

func main() {
	canvas := screen.NewCanvas(10, 5, '.')
}
```

Nice, we now have a canvas!
But we can't see it on screen. For that we need a **Render** function

---

## Render

In the context of our canvas, a render function is responsible for displaying the canvas on the screen.

- Render = take data/state -> draw pixels (or shapes, text, colors) on the screen
(in our case, text characters)

### Let's implement our simple Render function

We need to loop over the canvas width and height. For every character (all dots, for now), we print it to the screen. After each row, we jump to the next line.

The position of each character is defined by:
* the column index (x)
* the row index (y)

I’ll call them _xc_ and _yc_ (“x coordinate” and “y coordinate”).

```Go
func (c *canvas) Render() {
	for yc := 0; yc < c.Height; yc++ { // Y axis (rows)
		for xc := 0; xc < c.Width; xc++ { // X axis (columns)
			fmt.Print(string(c.Background)) // draw the background character
		}
		fmt.Println() // move to the next line after each row
	}
}
```

### Rendering the Canvas

```Go
package main

import "github.com/nellfs/go2d/internal/screen"

func main() {
	canvas := screen.NewCanvas(10, 5, '.')

	canvas.Render()
}
```

If we run this program, we can finally see the canvas in our terminal emulator:

```bash
nellfs@desktop:~/work/go2d> go run cmd/main.go
..........
..........
..........
..........
..........
nellfs@desktop:~/work/go2d>
```

-**_YAAYYYYY! WE DID IT!!_**

---

## Wrapping up

We’ve created a canvas and rendered it on the screen.
In the next part, we’ll learn how to draw shapes on this canvas.
