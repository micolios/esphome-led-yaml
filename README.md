# ESPHome Addressable LED YAML Generator

Create static effects for ESPHome addressable LED strips, matrices and segmented displays.

The generator runs entirely in your browser and produces ready-to-paste `addressable_lambda` YAML for ESPHome.

## Features

- Create static effects for a single addressable LED strip.
- Choose an individual colour for every LED.
- Create coloured segments, with an editable LED count and colour for each segment.
- Automatically divide a strip into evenly sized coloured segments.
- Create square or rectangular LED matrices.
- Configure row-major or column-major data direction.
- Support serpentine / zig-zag wiring for both rows and columns.
- Choose the starting corner of the LED matrix.
- Click or click-and-drag to select and deselect individual LEDs.
- Apply a selected colour to one LED, a group of LEDs, or every LED.
- See a live preview showing each LED’s physical strip index.
- Generate ready-to-use ESPHome `addressable_lambda` YAML.
- Copy the generated YAML to the clipboard with one click.

## Modes

### Single strip

Use this mode for a normal, non-segmented addressable LED strip.

1. Select **Single strip**.
2. Enter the total number of LEDs.
3. Click an LED to select it, or click and drag to select several.
4. Choose a colour and select **Apply to selection**.
5. Copy the generated YAML into your ESPHome configuration.

You can also use **Apply to all** to set every LED to the same colour.

### Segmented strip

Use this mode to create repeating colour bands, flags, indicators or simple patterns.

1. Select **Segmented strip**.
2. Enter the physical strip length.
3. Enter the number of divisions you want.
4. Select **Divide evenly**.
5. Adjust each segment’s colour or LED count if required.

The generator automatically creates an `it.range()` instruction for each segment, keeping the generated YAML compact.

For example, a 21-LED strip divided into seven segments produces:

```yaml
it.range(0, 3) = Color(255, 0, 0);
it.range(3, 6) = Color(255, 127, 0);
it.range(6, 9) = Color(255, 255, 0);
it.range(9, 12) = Color(0, 255, 0);
it.range(12, 15) = Color(0, 255, 255);
it.range(15, 18) = Color(0, 0, 255);
it.range(18, 21) = Color(139, 0, 255);
```

If the strip length does not divide exactly, the remaining LEDs are distributed across the earliest segments. For example, 22 LEDs divided into seven segments creates segment sizes of:

```text
4, 3, 3, 3, 3, 3, 3
```

### Square / rectangle

Use this mode for LED matrices, panels and grids.

Configure:

- **Width** — number of LEDs across the matrix.
- **Height** — number of LEDs down the matrix.
- **Data direction** — whether the strip is wired row by row or column by column.
- **Serpentine / zig-zag** — enable this if alternate rows or columns reverse direction.
- **Starting corner** — where LED 0 is physically located.

The preview shows the physical strip index assigned to every matrix cell, making it easier to check that the wiring direction matches your actual display.

For example, a 5 × 2 matrix using serpentine rows produces:

```text
0  1  2  3  4
9  8  7  6  5
```

The same matrix using serpentine columns produces:

```text
0  3  4  7  8
1  2  5  6  9
```

## Colour selection

For single-strip and matrix modes, you can:

- Click an LED to select or deselect it.
- Click and drag to select or deselect a run of LEDs.
- Use the colour picker or enter a hex value such as `#91cefa`.
- Apply the chosen colour to the current selection.
- Apply the chosen colour to every LED.
- Clear the current selection.
- Reset every LED to black.

The LED preview shows the physical strip index inside each LED, helping you identify the correct LED before applying a colour.

## Generated YAML

The tool generates a complete ESPHome `effects:` block using `addressable_lambda`.

For individual LEDs, it generates lines such as:

```yaml
it[0] = Color(91, 206, 250);
it[1] = Color(245, 169, 184);[1]
it[2] = Color(255, 255, 255);
```

For segmented strips, it uses ranges:

```yaml
it.range(0, 3) = Color(91, 206, 250);
it.range(3, 5) = Color(245, 169, 184);
```

Copy the generated YAML and paste it into the `effects:` section of an addressable ESPHome light configuration.

## Example ESPHome usage

```yaml
light:
  - platform: neopixelbus
    id: led_strip
    name: "LED Strip"
    pin: GPIO13
    num_leds: 21
    variant: WS2812

    effects:
      - addressable_lambda:
          name: "Custom Static"
          update_interval: 1s
          lambda: |-
            it.range(0, 3) = Color(255, 0, 0);
            it.range(3, 6) = Color(255, 127, 0);
            it.range(6, 9) = Color(255, 255, 0);
            it.range(9, 12) = Color(0, 255, 0);
            it.range(12, 15) = Color(0, 255, 255);
            it.range(15, 18) = Color(0, 0, 255);
            it.range(18, 21) = Color(139, 0, 255);
```

`update_interval: 1s` is suitable for a static effect. The lambda is executed when the effect starts and then once per update interval.

## Running locally

No build step is required.

1. Download or clone this repository.
2. Open `index.html` in a web browser.

## ESPHome compatibility

This generator is intended for ESPHome addressable lights, including:

- `neopixelbus`
- `wled`
- Other addressable light platforms that support `addressable_lambda`

The generated YAML uses ESPHome’s `addressable_lambda` effect, `it[index]`, and `it.range(start, end)`.
