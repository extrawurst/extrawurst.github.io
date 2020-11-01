---
title:  "Benchmark Godot & Rust"
date:   2020-11-01 17:00:00
categories: [general,gamedev,rust]
---

# The Contest

* drawing tons of lines

![res]({{ site.url }}/assets/godot-rust-benchmark/result.png)

# The Contenders

## GDScript

```gd
extends CanvasItem

export var cnt = 4000
export var start = Vector2(250,250)
export var rad = 200

func _draw():
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

![vs]({{ site.url }}/assets/godot-rust-benchmark/visualscript.png)

## GDNative (Rust)

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

yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada yada 