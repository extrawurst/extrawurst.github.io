---
title:  "Benchmark Godot & Rust"
date:   2020-11-07 18:00:00
categories: [general,gamedev,rust]
---

I recently started looking into using Rust and the [Godot](https://godotengine.org) Game Engine for developing games. A very quick experiment recently was to compare performance of GDScript, Visual scripting and Rust for the same task.

# The Contest

It is simple: Let us just draw a ton of lines to have a lot of traffic between our code and the engine:

![res]({{ site.url }}/assets/godot-rust-benchmark/result.png)

In fact it is so many lines that we end up with a filled circle. Nothing beautiful, this is just a benchmark afterall 👌

# The Contenders

Let us look at the contenders:
* gdscript
* visual script
* gdnative (rust)

**GDScript** is the official scripting language shipping with Godot.

**Visual Script** is the official node based way to visually script in Godot (think Unreal Blueprint or Unity Bolt)

**GDNative** is the official C-Api to access Godot from escentially any language. We are using [Godot-Rust](https://github.com/godot-rust/godot-rust) to be able to compile shared libraries from Rust code to interface with Godot.

---

The entire test with all three options can be found on github: [extrawurst/godot-rust-benchmark](https://github.com/extrawurst/godot-rust-benchmark)

Rerun it for yourself :)

## GDScript

Let's start with the official way of doing things in Godot - using GDScript:

```gd
extends CanvasItem

# how many lines to draw? (this can be adjusted from the editor UI)
export var cnt = 6000
# this is the center point
export var start = Vector2(250,250)
# radius (line length)
export var rad = 200

func _draw():
	# lets measure the runtime
	var startTime = OS.get_ticks_usec()

	var cntf = float(cnt)
	for n in range(cnt):
		var x = sin(n/cntf * 360.0)*rad
		var y = cos(n/cntf * 360.0)*rad
		draw_line(
			start, 
			start+Vector2(x, y), 
			Color(255, 0, 0), 
			1,
			false)
	
	print("bench: " + String(OS.get_ticks_usec() - startTime))

func _process(_delta):
	update()
```

## Visual Script

The following screenshot shows the same logic in a visual node based way:

![vs]({{ site.url }}/assets/godot-rust-benchmark/visualscript.png)

We immediately see how this is more verbose but at least it is possible and it even just crashed once on me 🙈

## GDNative (Rust)

We are using [Godot-Rust](https://github.com/godot-rust/godot-rust) for this.

```rust
#[export]
fn _draw(&self, owner: &CanvasItem) {
	let start_time = OS::godot_singleton().get_ticks_usec();

	let cntf = self.cnt as f32;

	for n in 0..self.cnt {
		let x = f32::sin(n as f32 / cntf * 360.0) * self.rad;
		let y = f32::cos(n as f32 / cntf * 360.0) * self.rad;
		let target = Vector2::new(x, y) + self.start;

		owner.draw_line(
			self.start, 
			target, 
			Color::rgb(0.0, 0.0, 1.0), 
			1.0, 
			false)
	}

	godot_print!(
		"bench: {}",
		OS::godot_singleton().get_ticks_usec() - start_time
	);
}
```

# The Results

I am not going to further comment on the ergonomics of either language. I really did this for two reasons: 1) can we do all we need in the visual script and 2) how does performance compare between the alternatives

Here are the timings:

| type | usecs | slowdown | 
|---|---|---|
| gdnative (rust) | ~1000 usec | - |
| gdscript | ~5000 usec | 5x |
| visual script | ~7000 usec | 7x |

<sub><sup>(executed on a macbook 2016 3,3 GHz i7, 16 GB Ram, Intel Iris 550 and Godot 3.2.3)</sup></sub>

On twitter people noted that this might change with Godot 4.0 and the support of type checking in gdscript. Thid could be interesting to measure once 4.0 is released. 

For now my conclusion is:

* GDScript is easy and quick to learn
* Visual Scripting in Godot works although feels a little instable
* Godot-Rust is a clear alternative to write entire Godot games in

Of course point 3) is limited to people coming with a Rust background otherwise the Rust part in it is a clear challenge to learn first.