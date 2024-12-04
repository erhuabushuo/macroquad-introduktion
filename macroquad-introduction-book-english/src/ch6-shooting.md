# Bullet hell

<div class="noprint">

![Screenshot](images/screenshots-web/shooting.gif#center)

</div>
<div class="onlyprint">

![Screenshot](images/screenshots-print/shooting.png#center)

</div>

It is slightly unfair that our poor circle isn't able to defend itself against
the terrifying squares. So it's time to implement the ability for the circle to
shoot bullets.

## Implementation

### Dead or alive?

To keep track of which squares have been hit by bullets we add the field
`collided` of the type `bool` to the struct `Shape`.

```rust [hl,6]
{{#include ../../my-game/examples/shooting.rs:shape}}
```

### Keeping track

We need another vector to keep track of all the bullets. For simplicity's sake
we'll call it `bullets`. Add it after the `squares` vector. Here we'll also
set the type of the elements to ensure that the Rust compiler knows what type
it is before we have added anything to it. We'll use the struct `Shape` for
the bullets as well.

```rust
{{#include ../../my-game/examples/shooting.rs:bullets}}
```

### Shoot bullets

After the circle has moved we'll add a check if the player has pressed the
space key and add a bullet to the `bullets` vector. The `x` and `y`
coordinates of the bullet are set to the same values as for the circle, and
the speed is set to twice that of the circle.

```rust
{{#include ../../my-game/examples/shooting.rs:shoot}}
```

```admonish note title="Only one shot"
Note that we're using the function `is_key_pressed()` which only returns true
during the frame when the key was first pressed.
```

Since we added a new field to the `Shape` struct we'll need to set it when we
create a square.

```rust [hl,6]
{{#include ../../my-game/examples/shooting.rs:squarecollided}}
```

### Move bullets

We don't want the bullets to be stationary mines, so we'll have to loop over
the `bullets` vector and move them in the `y` direction. Add the following
code after the code that moves the squares.

```rust [hl,4-6]
{{#include ../../my-game/examples/shooting.rs:movebullets}}
```

### Remove bullets and squares

Make sure to remove the bullets that have exited the screen in the same way
that the squares are removed.

```rust
{{#include ../../my-game/examples/shooting.rs:removebullets}}
```

Now it is time to remove all the squares and bullets that have collided. It
can be done with the `retain` method on the vectors which takes a predicate
that should return `true` if the element should be kept. We'll just check
whether the `collided` field on the struct is false. Do the same thing for
both the `squares` and the `bullets` vectors.

```rust
{{#include ../../my-game/examples/shooting.rs:removecollided}}
```

### Collision

After the check if the circle has collided with a square we'll add another
check if any of the squares have been hit by a bullet. We'll set the field
`collided` to true for both the square and the bullet so that they can be
removed.

```rust
{{#include ../../my-game/examples/shooting.rs:collided}}
```

### Clear bullets

When the game is over we also have to clear the `bullets` vector so that all
the bullets are removed when a new game is started.

```rust [hl,3]
{{#include ../../my-game/examples/shooting.rs:clearbullets}}
```

### Draw bullets

Before the circle is drawn we'll draw all the bullets that the player has
shot. This ensures that they are drawn behind all the other shapes.

```rust
{{#include ../../my-game/examples/shooting.rs:drawbullets}}
```

```admonish info title="Outlined circle"
The is another function called
[`draw_circle_lines()`](https://docs.rs/macroquad/latest/macroquad/shapes/fn.draw_circle_lines.html)
that can be used to draw a circle with just the outline.
```

This is all the code that is needed for the circle to be able to shoot down
all the fearsome squares.

```admonish tip title="Challenge: Bullet reloading" class="challenge"
To increase the difficulty it's possible to add a minimum time for reloading
between each shot. Try using the function
[`get_time()`](https://docs.rs/macroquad/latest/macroquad/time/fn.get_time.html)
to save when the last shot was fired and compare it with the current time.
Only add a bullet if the difference is above a certain threshold.

Another possibility is to only allow a specific number of bullets on the
screen at the same time.
```

<div class="noprint">

## Full source code

<details>
  <summary>Click to show the the full source code</summary>

```rust
{{#include ../../my-game/examples/shooting.rs:all}}
```
</details>
</div>

<div class="noprint">

## Quiz

Try your knowledge by answering the following quiz before you move on to the
next chapter.

{{#quiz ../quizzes/shooting.toml}}

</div>
